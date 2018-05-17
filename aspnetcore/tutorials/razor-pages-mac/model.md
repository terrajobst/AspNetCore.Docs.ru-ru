---
title: Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio для Mac
author: rick-anderson
description: Узнайте, как добавить модель в приложение Razor Pages в ASP.NET Core с помощью Visual Studio для Mac.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 97bc9f14b8d6da958a7f587e54a37d2d0e0aabd4
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a>Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio для Mac

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Добавление модели данных

* В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши, выберите пункт **Добавить** > **Новая папка**. Присвойте папке имя *Models*.
* Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить** > **Новый файл**.
* В диалоговом окне **Новый файл** выполните следующие действия.

  * Выберите на левой панели пункт **Общие**.
  * Выберите в центральной области **Пустой класс**.
  * Назовите класс **Movie** и выберите **Создать**.

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

Щелкните правой кнопкой мыши волнистую красную линию, например `MovieContext` в строке `services.AddDbContext<MovieContext>(options =>`. Выберите **Быстрое исправление > С помощью RazorPagesMovie.Models;**. Visual Studio добавляет с оператор using.

Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.

![Страница "Создать"](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Пакеты Entity Framework Core NuGet для миграций

Средства EF для интерфейса командной строки (CLI) доступны в [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Щелкните ссылку [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet), чтобы получить номер версии. Чтобы установить этот пакет, добавьте его в коллекцию `DotNetCliToolReference` в файле *.csproj*. **Примечание**. Необходимо установить этот пакет, отредактировав файл *.csproj*; использовать команду `install-package` или графический пользовательский интерфейс диспетчера пакетов нельзя.

Чтобы изменить файл *.csproj*, выполните следующие действия.

* Выберите **Файл** > **Открыть**, а затем выберите файл *.csproj*.
* Выберите **Параметры**.
* Изменение значение параметра **Открыть с помощью** на **Редактор исходного кода**.

![Изменение файла csproj](model/csproj.png)

Добавьте ссылку на инструмент `Microsoft.EntityFrameworkCore.Tools.DotNet` во вторую группу **\<ItemGroup>**.

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

Номера версий, показанные в приведенном ниже коде, были правильными на момент написания статьи.

[!INCLUDE [model3](../../includes/RP/model3.md)]

[!INCLUDE [model 4x](../../includes/RP/model4x.md)]

[!INCLUDE [model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE [model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a>Добавление файлов страниц и фильмов в проект

* В Visual Studio щелкните папку *Pages* правой кнопкой мыши и выберите пункт **Добавить > Добавить существующую папку**.
* Выберите папку *Movies*.
* В диалоговом окне *Выбор файлов для включения в проект* выберите **Включить все**.

В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.

> [!div class="step-by-step"]
> [Предыдущая статья — "Начало работы"](xref:tutorials/razor-pages-mac/razor-pages-start)
> [Следующая статья — "Сформированные страницы Razor Pages"](xref:tutorials/razor-pages-mac/page)
