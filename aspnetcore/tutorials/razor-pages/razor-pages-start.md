---
title: Учебник. Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
description: В этой серии руководств объясняется, как использовать Razor Pages в ASP.NET Core. Узнайте, как создать модель, сгенерировать код для Razor Pages, использовать Entity Framework Core и SQL Server для доступа к данным, добавлять функции поиска и проверки ввода, а также использовать возможность миграции для обновления модели.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 88449a0064dad42d8d2bf9fbdd67078e4c2ba8de
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210071"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>Учебник. Начало работы с Razor Pages в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Это первый учебник из серии. В этой [серии](xref:tutorials/razor-pages/index) приводятся основные сведения о сборке веб-приложения Razor Pages в ASP.NET Core.

[!INCLUDE[](~/includes/advancedRP.md)]

В конце серии вы получите приложение, которое управляет базой данных фильмов.  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

В этом учебнике рассмотрены следующие задачи.

> [!div class="checklist"]
> * Создание веб-приложения Razor Pages.
> * Запустите приложение.
> * Анализ файлов проекта.

В конце этого учебника вы получите рабочее веб-приложения Razor Pages, сборку которого вы будете выполнять в последующих учебниках.

![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a>Создание веб-приложения Razor Pages

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.

* Создайте новое веб-приложение ASP.NET Core. Назовите проект **RazorPagesMovie**. Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* Выберите в раскрывающемся списке **ASP.NET Core 2.2**, а затем **Веб-приложение**.

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  Создается следующий начальный проект:

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

* Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).

* Смените каталог (`cd`) на папку, в которой будет содержаться проект.

* Выполните следующие команды:

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * Команда `dotnet new` создает новый проект Razor Pages в папке *RazorPagesMovie*.
  * Команда `code` открывает папку *RazorPagesMovie* в новом экземпляре Visual Studio Code.

  Появится диалоговое окно с предупреждением **В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?**

* Выберите ответ **Да**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

Из терминала выполните следующие команды:

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
```

Указанные выше команды используют [интерфейс командной строки .NET Core](/dotnet/core/tools/dotnet) для создания проекта Razor Pages.

## <a name="open-the-project"></a>Открытие проекта

В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *RazorPagesMovie.csproj*.

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a>Запуск приложения

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.

  [!INCLUDE[](~/includes/trustCertVS.md)]

  Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение. В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`. Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера. Localhost обслуживает только веб-запросы с локального компьютера. Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

* Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.

  [!INCLUDE[](~/includes/trustCertVSC.md)]

  Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`. В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`. Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера. Localhost обслуживает только веб-запросы с локального компьютера.
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

Выберите **Выполнить > Запуск без отладки**, чтобы запустить приложение. Visual Studio запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.

[!INCLUDE[](~/includes/trustCertMac.md)]

<!-- End of VS tabs -->

---

* На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.

  Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  На следующем рисунке показано приложение после принятия отслеживания:

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

## <a name="examine-the-project-files"></a>Анализ файлов проекта

Ниже приведен обзор основных папок и файлов проекта, с которыми вы будете работать в последующих учебниках.

### <a name="pages-folder"></a>Папка Pages

Содержит страницы Razor и вспомогательные файлы. Каждая страница Razor — это пара файлов.

* Файл *.cshtml*, содержащий HTML-разметку с кодом C# и синтаксисом Razor.
* Файл *. cshtml.cs*, содержащий код C#, который обрабатывает события страницы.

Имена вспомогательных файлов начинаются с символа подчеркивания. Например, файл *_Layout.cshtml* настраивает элементы пользовательского интерфейса, общие для всех страниц. Этот файл настраивает меню навигации в верхней части страницы и уведомление об авторских правах в нижней части страницы. Для получения дополнительной информации см. <xref:mvc/views/layout>.

### <a name="wwwroot-folder"></a>Папка wwwroot

Содержит статические файлы, такие как HTML-файлы, файлы JavaScript и CSS-файлы. Для получения дополнительной информации см. <xref:fundamentals/static-files>.

### <a name="appsettingsjson"></a>appSettings.json

Содержит данные конфигурации, например строки подключения. Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.

### <a name="programcs"></a>Program.cs

Содержит точку входа для программы. Для получения дополнительной информации см. <xref:fundamentals/host/web-host>.

### <a name="startupcs"></a>Startup.cs

Содержит код, который настраивает поведение приложения, например, требуется ли согласие для файлов cookie. Для получения дополнительной информации см. <xref:fundamentals/startup>.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Версия руководства на YouTube](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a>Следующие шаги

В этом учебнике рассмотрены следующие задачи.

> [!div class="checklist"]
> * Создание веб-приложения Razor Pages.
> * Запуск приложения.
> * Анализ файлов проекта.

Перейдите к следующему учебнику в серии:

> [!div class="step-by-step"]
> [Добавление модели](xref:tutorials/razor-pages/model)
