---
title: "Добавление модели в приложение Razor Pages с помощью Visual Studio для Mac"
author: rick-anderson
description: "Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio для Mac"
keywords: "ASP.NET Core,Razor Pages,Razor,MVC,модель"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 648ecd3a782fa489b727982ce5f7a2087539bf38
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a>Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio для Mac

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Добавление модели данных

* В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши, выберите пункт **Добавить** > **Новая папка**. Присвойте папке имя *Models*.
* Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить** > **Новый файл**.
* В диалоговом окне **Новый файл** выполните следующие действия.

  * Выберите на левой панели пункт **Общие**.
  * Выберите в центральной области **Пустой класс**.
  * Назовите класс **Movie** и выберите **Создать**.

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

Щелкните правой кнопкой мыши волнистую красную линию, например `MovieContext` в строке `services.AddDbContext<MovieContext>(options =>`. Выберите **Быстрое исправление > С помощью RazorPagesMovie.Models;**. Visual Studio добавляет с оператор using.

Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.

![Страница "Создать"](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Пакеты Entity Framework Core NuGet для миграций

Средства EF для интерфейса командной строки (CLI) доступны в [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Чтобы установить этот пакет, добавьте его в коллекцию `DotNetCliToolReference` в файле *.csproj*. **Примечание**. Необходимо установить этот пакет, отредактировав файл *.csproj*; использовать команду `install-package` или графический пользовательский интерфейс диспетчера пакетов нельзя.

Чтобы изменить файл *.csproj*, выполните следующие действия.

* Выберите **Файл** > **Открыть**, а затем выберите файл *.csproj*.
* Выберите **Параметры**.
* Изменение значение параметра **Открыть с помощью** на **Редактор исходного кода**.

![Изменение файла csproj](model/csproj.png)

Добавьте ссылку на инструмент `Microsoft.EntityFrameworkCore.Tools.DotNet` во вторую группу **\<ItemGroup>**:

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?range=12-16&highlight=4)]

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a>Добавление файлов страниц и фильмов в проект

* В Visual Studio щелкните папку *Pages* правой кнопкой мыши и выберите пункт **Добавить > Добавить существующую папку**.
* Выберите папку *Movies*.
* В диалоговом окне *Выберите файлы для добавления в проект* выберите параметр **Включить все**.

В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.

>[!div class="step-by-step"]
[Назад: Начало работы](xref:tutorials/razor-pages-mac/razor-pages-start)
[Далее: Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)
