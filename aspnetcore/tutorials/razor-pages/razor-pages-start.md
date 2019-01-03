---
title: Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: В этой серии руководств объясняется, как использовать Razor Pages в ASP.NET Core. Узнайте, как создать модель, сгенерировать код для Razor Pages, использовать Entity Framework Core и SQL Server для доступа к данным, добавлять функции поиска и проверки ввода, а также использовать возможность миграции для обновления модели.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1152ebfcee48a46ecd28c941fce32d3fc1e05c41
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861632"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>Руководство. Начало работы с Razor Pages в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом учебнике приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core.

Это приложение служит для управления базой данных названий фильмов. Вы научитесь:

> [!div class="checklist"]
> * Создание веб-приложения Razor Pages.
> * Добавление модели и формирование шаблона.
> * Работа с базой данных.
> * Добавление поиска и проверки.

В конечном итоге вы получите приложение, позволяющее управлять названиями фильмов и отображать их.

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-web-app"></a>Создание веб-приложения Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.
* Создайте новое веб-приложение ASP.NET Core. Назовите проект **RazorPagesMovie**. Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.
 ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* Выберите в раскрывающемся списке **ASP.NET Core 2.2**, а затем **Веб-приложение**.

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  Создается следующий начальный проект:

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

* Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.

  Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение. В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`. Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера. Localhost обслуживает только веб-запросы с локального компьютера. Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт. На представленном выше снимке экрана используется номер порта 5001. При запуске приложения вы увидите другой номер порта.

  Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде. Многие разработчики предпочитают использовать режим без отладки, чтобы быстро обновлять страницу и просматривать изменения.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

* Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Смените каталог (`cd`) на папку, в которой будет содержаться проект.
* Выполните следующую команду:

   ```console
   dotnet new webapp -o RazorPagesMovie
   code -r RazorPagesMovie
   ```

  * Появится диалоговое окно с предупреждением **В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?**  Выберите ответ **Да**.

  * `dotnet new webapp -o RazorPagesMovie`. Создает новый проект Razor Pages в папке *RazorPagesMovie*.
  * `code -r RazorPagesMovie`. Загружает файл проекта *RazorPagesMovie.csproj* в Visual Studio Code.

### <a name="launch-the-app"></a>Запуск приложения

* Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.

  Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`. В адресной строке указывается `localhost:port:5001`, а не что-либо типа `example.com`. Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера. Localhost обслуживает только веб-запросы с локального компьютера.

  Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде. Многие разработчики предпочитают использовать режим без отладки, чтобы быстро обновлять страницу и просматривать изменения.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

Из терминала выполните следующие команды:

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Указанные выше команды используют [интерфейс командной строки .NET Core](/dotnet/core/tools/dotnet) для создания и запуска проекта Razor Pages. Откройте в браузере страницу http://localhost:5000 для просмотра приложения.

## <a name="open-the-project"></a>Открытие проекта

Нажмите клавиши CTRL+C, чтобы завершить работу приложения.

В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *RazorPagesMovie.csproj*.

### <a name="launch-the-app"></a>Запуск приложения

Выберите **Выполнить > Запуск без отладки**, чтобы запустить приложение. Visual Studio запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.

<!-- End of VS tabs -->

---

* Нажмите **Принять**, чтобы согласиться на отслеживание. Это приложение не отслеживает персональные данные. Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  На следующем рисунке показано приложение после принятия отслеживания:

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

## <a name="project-files-and-folders"></a>Защита файлов и папок

В следующей таблице перечислены файлы и папки в проекте. На этой стадии важнее всего разобраться с файлом *Startup.cs*. Каждую из указанных ниже ссылок просматривать не требуется. Ссылки приводятся для справки и позволяют получить дополнительную информацию о каком-либо файле или папке в проекте.

| Файл или папка              | Цель |
| ----------------- | ------------ |
| *wwwroot* | Содержит статические файлы. См. [Статические файлы](xref:fundamentals/static-files). |
| *Pages* | Папка для [Razor Pages](xref:razor-pages/index). |
| *appsettings.json* | [Конфигурация](xref:fundamentals/configuration/index) |
| *Program.cs* | [Содержит](xref:fundamentals/host/index) приложение ASP.NET Core.|
| *Startup.cs* | Настраивает службы и конвейер обработки запросов. См. раздел [Запуск](xref:fundamentals/startup).|

### <a name="the-pages-folder"></a>Папка "Pages"

Файл *_Layout.cshtml* содержит общие элементы HTML (скрипты и таблицу стилей) и определяет макет для приложения. Например, при выборе ссылки **RazorPagesMovie**, **Главная** или **Конфиденциальность** отображаются одни и те же элементы. В число общих элементов входят меню навигации наверху экрана и заголовок внизу окна. Дополнительные сведения см. в статье о [макете](xref:mvc/views/layout).

Файл *_ViewImports.cshtml* содержит директивы Razor, импортированные в каждую страницу Razor Pages. Дополнительные сведения см. в разделе [Импорт общих директив](xref:mvc/views/layout#importing-shared-directives).

Файл *_ViewStart.cshtml* определяет свойство Razor Pages `Layout`, необходимое для использования файла *_Layout.cshtml*. Дополнительные сведения см. в статье о [макете](xref:mvc/views/layout).

Файл *_ValidationScriptsPartial.cshtml* содержит ссылку на сценарии проверки [jQuery](https://jquery.com/). Когда далее в этом учебнике мы добавим страницы `Create` и `Edit`, будет использоваться файл *_ValidationScriptsPartial.cshtml*.

Страницы `Index`, `Error` и `Privacy` имеют следующее назначение:

* `Index`. Запуск приложения.
* `Error`. Вывод сведений об ошибке.
* `Privacy`. Вывод сведений о политике конфиденциальности сайта.

В рамках этого учебника предыдущие страницы не используются.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

<a name="f7"></a>
### <a name="use-f7-to-toggle-between-a-razor-page-and-the-pagemodel"></a>Переключения между Razor Pages и PageModel с помощью клавиши F7

Клавиша F7 позволяет включить переключение между Razor Pages (файл *\*.cshtml*) и файлом C# (*\*.cshtml.cs*).

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

<!-- TODO review  Need something in these tabs -->

По соглашению Razor Page (файл *\*.cshtml*) и связанная `PageModel` имеют одинаковое имя корневого файла.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

По соглашению Razor Page (файл *\*.cshtml*) и связанная `PageModel` имеют одинаковое имя корневого файла.

---

> [!div class="step-by-step"]
> [Далее: добавление модели](xref:tutorials/razor-pages/model)